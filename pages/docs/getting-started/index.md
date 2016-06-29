---
title: Getting Started
---

## Prerequisites

Scorex requires Java 1.8. If you don’t have the JDK, you have to install it from [Oracle`s JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) Site.

## Modules
Scorex is very modular and consists of several JARs containing different features.
* scorex-basics - framework core.
* scorex-transaction - implementation of the transactional layer.
* scorex-consensus - Nxt and Quora consensus implementations.
* scorex-perma - Permacoin consensus implementation.

## Adding Scorex to your project

To include Scorex in an existing project use the library published on Maven Central. For sbt projects add required modules to your build definition - build.sbt or project/Build.scala:

```scala
libraryDependencies ++= Seq(
  "org.consensusresearch" %% "scorex-basics" % "1.+",
  "org.consensusresearch" %% "scorex-consensus" % "1.+",
  "org.consensusresearch" %% "scorex-perma" % "1.+",
  "org.consensusresearch" %% "scorex-transaction" % "1.+"
)
```

For Maven projects add required modules to your <dependencies> like:

```html
<dependency>
  <groupId>org.consensusresearch</groupId>
  <artifactId>scorex-basics_2.11</artifactId>
  <version>1.2.5</version>
</dependency>
```

**Nightly builds**

Unstable nightly Scorex snapshots are published to [Sonatype](https://oss.sonatype.org/content/repositories/snapshots/org/consensusresearch/). To use them add Sonatype snapshots repo to your resolvers.

```scala
resolvers += "SonaType snapshots" at "https://oss.sonatype.org/content/repositories/snapshots"
```

## Running Scorex

Compose your application with modules, provided by Scorex framework

```scala
import akka.actor.Props
import scorex.api.http._
import scorex.app.Application
import scorex.consensus.nxt.NxtLikeConsensusModule
import scorex.consensus.nxt.api.http.NxtConsensusApiRoute
import scorex.network._
import scorex.settings.Settings
import scorex.transaction._

import scala.reflect.runtime.universe._


class MyApplication(val settingsFilename: String) extends Application {

  // Your application config
  override val applicationName = "my cool application"
  override val appVersion = ApplicationVersion(0, 1, 0)
  override implicit lazy val settings = new Settings with TransactionSettings {
    override val filename: String = settingsFilename
  }

  // Define consensus and transaction modules of your application
  override implicit lazy val consensusModule = new NxtLikeConsensusModule()
  override implicit lazy val transactionModule= new SimpleTransactionModule()(settings, this)

  // Define API routes of your application
  override lazy val apiRoutes = Seq(
    BlocksApiRoute(this),
    TransactionsApiRoute(this),
    new NxtConsensusApiRoute(this),
    WalletApiRoute(this),
    PaymentApiRoute(this),
    UtilsApiRoute(this),
    PeersApiRoute(this),
    AddressApiRoute(this)
  )

  // API types are required for swagger support
  override lazy val apiTypes = Seq(
    typeOf[BlocksApiRoute],
    typeOf[TransactionsApiRoute],
    typeOf[NxtConsensusApiRoute],
    typeOf[WalletApiRoute],
    typeOf[PaymentApiRoute],
    typeOf[UtilsApiRoute],
    typeOf[PeersApiRoute],
    typeOf[AddressApiRoute]
  )

  // Create your custom messages and add them to additionalMessageSpecs
  override lazy val additionalMessageSpecs = TransactionalMessagesRepo.specs

  // Start additional actors
  actorSystem.actorOf(Props(classOf[UnconfirmedPoolSynchronizer], this))
}
```

Create json file with your [settings](/docs/settings/).

Run your application by simple runner

```scala
object MyApplication extends App {

  // Provide filename by command-line arguments
  val filename = args.headOption.getOrElse("settings.json")
  // Create application instance
  val application = new MyApplication(filename)
  // Run it
  application.run()
  // Generate account in your wallet
  if (application.wallet.privateKeyAccounts().isEmpty) application.wallet.generateNewAccounts(1)
}
```

Try it's API with swagger generated page, located at [http://host:port/swagger](http://host:port/swagger).
