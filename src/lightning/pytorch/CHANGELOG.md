Warning: truncated output (original token count: 127548)
Total output lines: 5017

# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/).

---

## [unreleased] - YYYY-MM-DD

### Added

- Added device, node, process, and training dataloader worker counts to full checkpoints as `training_metadata` ([#9123](https://github.com/Lightning-AI/pytorch-lightning/issues/9123))

- Added `suggest_integrations` flag to `Trainer` to control whether optional integration suggestions (e.g., litmodels, litlogger) are shown in logs ([#21632](https://github.com/Lightning-AI/pytorch-lightning/pull/21632))

- Fixed ``ModelParallelStrategy`` single-file checkpointing when ``torch.compile`` wraps the model so optimizer states no longer raise ``KeyError`` during save ([#21357](https://github.com/Lightning-AI/pytorch-lightning/issues/21357))

- Fixed gradient clipping not working with fused optimizers when using ``bf16-mixed`` precision ([#21435](https://github.com/Lightning-AI/pytorch-lightning/issues/21435))

- Added `log_key_prefix` parameter to `LearningRateMonitor` callback for prefixing logged metric names ([#21612](https://github.com/Lightning-AI/pytorch-lightning/issues/21612))

- Added `log_key_prefix` parameter to `Trainer` to control the prefix of Trainer-generated metric keys (e.g. `epoch`) ([#21782](https://github.com/Lightning-AI/pytorch-lightning/issues/21782))

- Added a `PossibleUserWarning` when the number of training batches is smaller than `accumulate_grad_batches` ([#21811](https://github.com/Lightning-AI/pytorch-lightning/pull/21811))

- Added support for remote storage (fsspec URLs) when saving and loading distributed checkpoints with `FSDPStrategy` ([#21775](https://github.com/Lightning-AI/pytorch-lightning/pull/21775))

- Added support for remote storage (fsspec URLs) when saving and loading distributed checkpoints with `ModelParallelStrategy` ([#21797](https://github.com/Lightning-AI/pytorch-lightning/issues/21797))

### Changed

- Changed the `dirpath` argument of `lightning.pytorch.profilers` profilers to accept `str | os.PathLike[str]` instead of `str | pathlib.Path` ([#21871](https://github.com/Lightning-AI/pytorch-lightning/pull/21871))

### Removed

- Removed dead code paths for PyTorch below 2.6 ([#21870](https://github.com/Lightning-AI/pytorch-lightning/pull/21870))

### Fixed

- Fixed `LightningCLI` emitting `jsonargparse` deprecation warnings ([#21900](https://github.com/Lightning-AI/pytorch-lightning/issues/21900))

- Fixed `RichProgressBar` showing a nonsensical negative epoch total (e.g. `Epoch 5/-2`) when `Trainer(max_epochs=-1)` (unlimited epochs) is used and training stops via another condition ([#21925](https://github.com/Lightning-AI/pytorch-lightning/issues/21925))

- Fixed `FSDPPrecision` rejecting a user-provided gradient scaler with `16-mixed` precision, and dropping it instead of keeping it ([#21831](https://github.com/Lightning-AI/pytorch-lightning/pull/21831))

- Fixed crash when calling ``self.log()`` inside a ``torch.compile``-wrapped ``LightningModule`` on PyTorch 2.12/2.13 by disabling Dynamo tracing at the ``LightningModule.log`` boundary ([#21836](https://github.com/Lightning-AI/pytorch-lightning/issues/21836))

- Fixed PyTorch Lightning profiler not capturing dataloader worker initialization time ([#21771](https://github.com/Lightning-AI/pytorch-lightning/issues/21771))

- Fixed `FSDPStrategy` raising `RuntimeError` under PyTorch 2.5+ when `root_device` is CPU, by passing an explicit `torch.device("cpu")` instead of `device_id=None` (relevant only when the GPU-accelerator guard is bypassed) ([#21774](https://github.com/Lightning-AI/pytorch-lightning/pull/21774))

- Fixed non-zero process exits in `CombinedLoader.reset()` with large tensors and persistent spawned workers by avoiding explicit `_shutdown_workers()` calls and relying on iterator cleanup via `del` [#21708](https://github.com/Lightning-AI/pytorch-lightning/issues/21708)

- Fixed `SIGTERMException` producing a zero exit code instead of 143 (128 + SIGTERM) ([#21623](https://github.com/Lightning-AI/pytorch-lightning/issues/21623))

- fixed AccumulateGrad stream mismatch warning when using DDP with Trainer ([#21746](https://github.com/Lightning-AI/pytorch-lightning/pull/21746))

- Fixed `LightningModule.toggle_optimizer` / `untoggle_optimizer` breaking under `torch.compile` by disabling Dynamo tracing on these bookkeeping helpers ([#21513](https://github.com/Lightning-AI/pytorch-lightning/issues/21513))

- Fixed `CUDAAccelerator.setup_device` initializing CUDA on an unrelated device by calling `torch.cuda.set_device` before the matmul precision check ([#21726](https://github.com/Lightning-AI/pytorch-lightning/pull/21726))

---

## [2.6.6] - 2026-09-10

### Fixed

- Fixed arbitrary code execution in `load_from_checkpoint` by restricting the `_instantiator` hyperparameter to an allowlist of trusted instantiators ([#21832](https://github.com/Lightning-AI/pytorch-lightning/pull/21832))
- Fixed arbitrary code execution in `load_from_checkpoint` by rejecting a checkpoint `_class_path` that does not resolve to an already imported subclass of the loaded class ([#21914](https://github.com/Lightning-AI/pytorch-lightning/pull/21914))

---

## [2.6.4] - 2026-05-20

> Versions 2.6.2 and 2.6.3 were skipped due to a supply chain security compromise. See [#21691](https://github.com/Lightning-AI/pytorch-lightning/issues/21691) for details.

### Removed

- Removed support for Neptune logger ([#21572](https://github.com/Lightning-AI/pytorch-lightning/pull/21572))

### Changed

- Raise minimum `LitLogger` version to `2026-03-17` ([#21591](https://github.com/Lightning-AI/pytorch-lightning/pull/21591))

### Fixed

- Fixed `val_check_interval` raising `ValueError` when `limit_val_batches=0` and interval exceeds training batches ([#21560](https://github.com/Lightning-AI/pytorch-lightning/pull/21560))
- Fixed pkg-resources deprecation issue ([#21538](https://github.com/Lightning-AI/pytorch-lightning/pull/21538))
- Fixed FSDP mixed precision (`bf16-mixed`, `16-mixed`) initializing model parameters in half precision instead of fp32 ([#21586](https://github.com/Lightning-AI/pytorch-lightning/pull/21586))
- Fixed `device_mesh` type hint in `FSDPStrategy` to accept a 2-element tuple via the CLI ([#21581](https://github.com/Lightning-AI/pytorch-lightning/pull/21581))
- Fixed `RichModelSummary` model size display formatting ([#21467](https://github.com/Lightning-AI/pytorch-lightning/pull/21467))
- Fixed floating-point precision in `SimpleProfiler` duration aggregation by using `math.fsum` ([#21525](https://github.com/Lightning-AI/pytorch-lightning/pull/21525))

---

## [2.6.1] - 2026-01-30

### Added

- Added method chaining support to `LightningModule.freeze()` and `LightningModule.unfreeze()` by returning `self` ([#21469](https://github.com/Lightning-AI/pytorch-lightning/pull/21469))
- Added litlogger integration([#21430](https://github.com/Lightning-AI/pytorch-lightning/pull/21430))

### Deprecated

- Deprecated `to_torchscript` method due to deprecation of TorchScript in PyTorch ([#21397](https://github.com/Lightning-AI/pytorch-lightning/pull/21397))


### Removed

- Removed support for Python 3.9 due to end-of-life status ([#21398](https://github.com/Lightning-AI/pytorch-lightning/pull/21398))

### Fixed

- Fixed `save_hyperparameters(ignore=...)` behavior so subclass ignore rules override base class rules (#[21490](https://github.com/Lightning-AI/pytorch-lightning/pull/21490))
- Fixed `LightningDataModule.load_from_checkpoint` to restore the datamodule subclass and hyperparameters ([#21478](https://github.com/Lightning-AI/pytorch-lightning/pull/21478))
- Fixed ``ModelParallelStrategy`` single-file checkpointing when ``torch.compile`` wraps the model so optimizer states no longer raise ``KeyError`` during save ([#21357](https://github.com/Lightning-AI/pytorch-lightning/issues/21357))
- Sanitize profiler filenames when saving to avoid crashes due to invalid characters ([#21395](https://github.com/Lightning-AI/pytorch-lightning/pull/21395))
- Fixed `StochasticWeightAveraging` with infinite epochs ([#21396](https://github.com/Lightning-AI/pytorch-lightning/pull/21396))
- Fixed `_generate_seed_sequence_sampling` function not producing unique seeds ([#21399](https://github.com/Lightning-AI/pytorch-lightning/pull/21399))
- Fixed `ThroughputMonitor` callback emitting warnings too frequently ([#21453](https://github.com/Lightning-AI/pytorch-lightning/pull/21453))


## [2.6.0] - 2025-11-28

### Added

- Added `WeightAveraging` callback that wraps the PyTorch `AveragedModel` class ([#20545](https://github.com/Lightning-AI/pytorch-lightning/pull/20545))
- Added Torch-Tensorrt integration with `LightningModule` ([#20808](https://github.com/Lightning-AI/pytorch-lightning/pull/20808))
- Added time-based validation support though `val_check_interval` ([#21071](https://github.com/Lightning-AI/pytorch-lightning/pull/21071))
- Added attributes to access stopping reason in `EarlyStopping` callback ([#21188](https://github.com/Lightning-AI/pytorch-lightning/pull/21188))
- Added support for variable batch size in `ThroughputMonitor` ([#20236](https://github.com/Lightning-AI/pytorch-lightning/pull/20236))
- Added `EMAWeightAveraging` callback that wraps Lightning's `WeightAveraging` class ([#21260](https://github.com/Lightning-AI/pytorch-lightning/pull/21260))

### Changed

- Expose `weights_only` argument for `Trainer.{fit,validate,test,predict}` and let `torch` handle default value ([#21072](https://github.com/Lightning-AI/pytorch-lightning/pull/21072))
- Default to `RichProgressBar` and `RichModelSummary` if the rich package is available. Fallback to TQDMProgressBar and ModelSummary otherwise ([#20896](https://github.com/Lightning-AI/pytorch-lightning/pull/20896))
- Add MPS accelerator support for mixed precision ([#21209](https://github.com/Lightning-AI/pytorch-lightning/pull/21209))

### Fixed

- Fixed edgecase when `max_trials` is reached in `Tuner.scale_batch_size` ([#21187](https://github.com/Lightning-AI/pytorch-lightning/pull/21187))
- Fixed case where `LightningCLI` could not be initialized with `trainer_default` containing callbacks ([#21192](https://github.com/Lightning-AI/pytorch-lightning/pull/21192))
- Fixed missing reset when `ModelPruning` is applied with lottery ticket hypothesis ([#21191](https://github.com/Lightning-AI/pytorch-lightning/pull/21191))
- Fixed preventing recursive symlink creation iwhen `save_last='link'` and `save_top_k=-1` ([#21186](https://github.com/Lightning-AI/pytorch-lightning/pull/21186))
- Fixed `last.ckpt` being created and not linked to another checkpoint ([#21244](https://github.com/Lightning-AI/pytorch-lightning/pull/21244))
- Fixed bug that prevented `BackboneFinetuning` from being used together with `LearningRateFinder` ([#21224](https://github.com/Lightning-AI/pytorch-lightning/pull/21224))
- Fixed `ModelPruning` sparsity logging bug that caused incorrect sparsity percentages ([#21223](https://github.com/Lightning-AI/pytorch-lightning/pull/21223))
- Fixed `LightningCLI` loading of hyperparameters from `ckpt_path` failing for subclass model mode ([#21246](https://github.com/Lightning-AI/pytorch-lightning/pull/21246))
- Fixed check the init args only when the given frames are in `__init__` method ([#21227](https://github.com/Lightning-AI/pytorch-lightning/pull/21227))
- Fixed how `ThroughputMonitor` calculated training time ([#21291](https://github.com/Lightning-AI/pytorch-lightning/pull/21291))
- Fixed synchronization of gradients in manual optimization with `DDPStrategy(static_graph=True)` ([#21251](https://github.com/Lightning-AI/pytorch-lightning/pull/21251))
- Fixed FSDP mixed precision semantics and added user warning ([#21361](https://github.com/Lightning-AI/pytorch-lightning/pull/21361))
- Fixed `ModelCheckpoint.file_exists` using broadcast in DDP, reducing memory usage when checking for existing checkpoints ([#19674](https://github.com/Lightning-AI/pytorch-lightning/issues/19674))


## [2.5.6] - 2025-11-05

### Changed

- Add `name()` function to accelerator interface (([#21325](https://github.com/Lightning-AI/pytorch-lightning/pull/21325)))

### Removed

- Remove support for deprecated and archived lightning-habana package ([#21327](https://github.com/Lightning-AI/pytorch-lightning/pull/21327))


## [2.5.5] - 2025-09-05

### Changed

- Include `exclude_frozen_parameters` to `DeepSpeedStrategy` ([#21060](https://github.com/Lightning-AI/pytorch-lightning/pull/21060))
- Include `PossibleUserWarning` that is raised if modules are in eval mode when training starts ([#21146](https://github.com/Lightning-AI/pytorch-lightning/pull/21146))

### Fixed

- Fixed `LightningCLI` not using `ckpt_path` hyperparameters to instantiate classes ([#21116](https://github.com/Lightning-AI/pytorch-lightning/pull/21116))
- Fixed callbacks by defer step/time-triggered `ModelCheckpoint` saves until validation metrics are available ([#21106](https://github.com/Lightning-AI/pytorch-lightning/pull/21106))
- Fixed with adding a missing device id for pytorch 2.8 ([#21105](https://github.com/Lightning-AI/pytorch-lightning/pull/21105))
- Fixed `TQDMProgressBar` not resetting correctly when using both a finite and iterable dataloader ([#21147](https://github.com/Lightning-AI/pytorch-lightning/pull/21147))
- Fixed cleanup of temporary files from `Tuner` on crashes ([#21162](https://github.com/Lightning-AI/pytorch-lightning/pull/21162))


## [2.5.4] - 2025-08-29

### Fixed

- Fixed `AsyncCheckpointIO` snapshots tensors to avoid race with parameter mutation ([#21079](https://github.com/Lightning-AI/pytorch-lightning/pull/21079))
- Fixed `AsyncCheckpointIO` threadpool exception if calling fit or validate more than one ([#20952](https://github.com/Lightning-AI/pytorch-lightning/pull/20952))
- Fixed learning rate not being correctly set after using `LearningRateFinder` callback ([#21068](https://github.com/Lightning-AI/pytorch-lightning/pull/21068))
- Fixed misalignment column while using rich model summary in `DeepSpeedstrategy` ([#21100](https://github.com/Lightning-AI/pytorch-lightning/pull/21100))
- Fixed `RichProgressBar` crashing when sanity checking using val dataloader with 0 len ([#21108](https://github.com/Lightning-AI/pytorch-lightning/pull/21108))

## [2.5.3] - 2025-08-13

### Changed

- Added `save_on_exception` option to `ModelCheckpoint` Callback ([#20916](https://github.com/Lightning-AI/pytorch-lightning/pull/20916))
- Allow `dataloader_idx_` in log names when `add_dataloader_idx=False` ([#20987](https://github.com/Lightning-AI/pytorch-lightning/pull/20987))
- Allow returning `ONNXProgram` when calling `to_onnx(dynamo=True)` ([#20811](https://github.com/Lightning-AI/pytorch-lightning/pull/20811))
- Extended support for general mappings being returned from `training_step` when using manual optimization ([#21011](https://github.com/Lightning-AI/pytorch-lightning/pull/21011))

### Fixed

- Fixed Allowing trainer to accept CUDAAccelerator instance as accelerator with FSDP strategy ([#20964](https://github.com/Lightning-AI/pytorch-lightning/pull/20964))
- Fixed progress bar console clearing for Rich `14.1+` ([#21016](https://github.com/Lightning-AI/pytorch-lightning/pull/21016))
- Fixed `AdvancedProfiler` to handle nested profiling actions for Python 3.12+ ([#20809](https://github.com/Lightning-AI/pytorch-lightning/pull/20809))
- Fixed rich progress bar error when resume training ([#21000](https://github.com/Lightning-AI/pytorch-lightning/pull/21000))
- Fixed double iteration bug when resumed from a checkpoint. ([#20775](https://github.com/Lightning-AI/pytorch-lightning/pull/20775))
- Fixed support for more dtypes in `ModelSummary` ([#21034](https://github.com/Lightning-AI/pytorch-lightning/pull/21034))
- Fixed metrics in `RichProgressBar` being updated according to user provided `refresh_rate` ([#21032](https://github.com/Lightning-AI/pytorch-lightning/pull/21032))
- Fixed `save_last` behavior in the absence of validation ([#20960](https://github.com/Lightning-AI/pytorch-lightning/pull/20960))
- Fixed integration between `LearningRateFinder` and `EarlyStopping` ([#21056](https://github.com/Lightning-AI/pytorch-lightning/pull/21056))
- Fixed gradient calculation in `lr_finder` for `mode="exponential"`  ([#21055](https://github.com/Lightning-AI/pytorch-lightning/pull/21055))
- Fixed `save_hyperparameters` crashing with `dataclasses` using `init=False` fields ([#21051](https://github.com/Lightning-AI/pytorch-lightning/pull/21051))


## [2.5.2] - 2025-06-20

### Changed

- Add `enable_autolog_hparams` argument to Trainer ([#20593](https://github.com/Lightning-AI/pytorch-lightning/pull/20593))
- Add `toggled_optimizer(optimizer)` method to the LightningModule, which is a context manager version of `toggle_optimize` and `untoggle_optimizer` ([#20771](https://github.com/Lightning-AI/pytorch-lightning/pull/20771))
- For cross-device local checkpoints, instruct users to install `fsspec>=2025.5.0` if unavailable ([#20780](https://github.com/Lightning-AI/pytorch-lightning/pull/20780))
- Check param is of `nn.Parameter` type for pruning sanitization ([#20783](https://github.com/Lightning-AI/pytorch-lightning/pull/20783))

### Fixed

- Fixed `save_hyperparameters` not working correctly with `LightningCLI` when there are parsing links applied on instantiation ([#20777](https://github.com/Lightning-AI/pytorch-lightning/pull/20777))
- Fixed `logger_connector` has an edge case where step can be a float ([#20692](https://github.com/Lightning-AI/pytorch-lightning/pull/20692))
- Fixed Synchronize SIGTERM Handling in DDP to Prevent Deadlocks ([#20825](https://github.com/Lightning-AI/pytorch-lightning/pull/20825))
- Fixed case-sensitive model name ([#20661](https://github.com/Lightning-AI/pytorch-lightning/pull/20661))
- CLI: resolve jsonargparse deprecation warning ([#20802](https://github.com/Lightning-AI/pytorch-lightning/pull/20802))
- Fix: move `check_inputs` to the target device if available during `to_torchscript` ([#20873](https://github.com/Lightning-AI/pytorch-lightning/pull/20873))
- Fixed progress bar display to correctly handle iterable dataset and `max_steps` during training ([#20869](https://github.com/Lightning-AI/pytorch-lightning/pull/20869))
- Fixed problem for silently supporting `jsonnet` ([#20899](https://github.com/Lightning-AI/pytorch-lightning/pull/20899))


## [2.5.1] - 2025-03-18

### Changed

- Allow LightningCLI to use a customized argument parser class ([#20596](https://github.com/Lightning-AI/pytorch-lightning/pull/20596))
- Change `wandb` default x-axis to `tensorboard`'s `global_step` when `sync_tensorboard=True` ([#20611](https://github.com/Lightning-AI/pytorch-lightning/pull/20611))
- CometML logger was updated to support the recent Comet SDK ([#20275](https://github.com/Lightning-AI/pytorch-lightning/pull/20275))
- bump: testing with latest `torch` 2.6 ([#20509](https://github.com/Lightning-AI/pytorch-lightning/pull/20509))

### Fixed

- Fixed CSVLogger logging hyperparameter at every write which increase latency  ([#20594](https://github.com/Lightning-AI/pytorch-lightning/pull/20594))
- Fixed OverflowError when resuming from checkpoint with an iterable dataset ([#20565](https://github.com/Lightning-AI/pytorch-lightning/issues/20565))
- Fixed swapped _R_co and _P to prevent type error ([#20508](https://github.com/Lightning-AI/pytorch-lightning/issues/20508))
- Always call `WandbLogger.experiment` first in `_call_setup_hook` to ensure `tensorboard` logs can sync to `wandb` ([#20610](https://github.com/Lightning-AI/pytorch-lightning/pull/20610))
- Fixed TBPTT example ([#20528](https://github.com/Lightning-AI/pytorch-lightning/pull/20528))
- Fixed test compatibility as AdamW became subclass of Adam ([#20574](https://github.com/Lightning-AI/pytorch-lightning/pull/20574))
- Fixed file extension of model checkpoints uploaded by NeptuneLogger ([#20581](https://github.com/Lightning-AI/pytorch-lightning/pull/20581))
- Reset trainer variable `should_stop` when `fit` is called ([#19177](https://github.com/Lightning-AI/pytorch-lightning/pull/19177))
- Fixed making `WandbLogger` upload models from all `ModelCheckpoint` callbacks, not just one ([#20191](https://github.com/Lightning-AI/pytorch-lightning/pull/20191))
- Error when logging to MLFlow deleted experiment ([#20556](https://github.com/Lightning-AI/pytorch-lightning/pull/20556))


## [2.5.0] - 2024-12-19

### Added

- Added `step` parameter to `TensorBoardLogger.log_hyperparams` to visualize changes during training ([#20176](https://github.com/Lightning-AI/pytorch-lightning/pull/20176))
- Added `str` method to datamodule ([#20301](https://github.com/Lightning-AI/pytorch-lightning/pull/20301))
- Added timeout to DeepSpeedStrategy ([#20474](https://github.com/Lightning-AI/pytorch-lightning/pull/20474))
- Added doc for Truncated Back-Propagation Through Time ([#20422](https://github.com/Lightning-AI/pytorch-lightning/pull/20422))
- Added FP8 + FSDP2 + torch.compile examples for PyTorch Lightning ([#20440](https://github.com/Lightning-AI/pytorch-lightning/pull/20440))
- Added profiling to `Trainer.save_checkpoint` ([#20405](https://github.com/Lightning-AI/pytorch-lightning/pull/20405))
- Added after_instantiate_classes hook to CLI ([#20401](https://github.com/Lightning-AI/pytorch-lightning/pull/20401))

### Changed

- Updated checkpointing documentation to mark `resume_from_checkpoint` as deprecated ([#20477](https://github.com/Lightning-AI/pytorch-lightning/pull/20477))
- Made plugin type checks more flexible ([#20186](https://github.com/Lightning-AI/pytorch-lightning/pull/20186))
- Changed seeding NumPy using `np.random.SeedSequence()` in `pl_worker_init_function()` to robustly seed NumPy-dependent dataloader workers ([#20369](https://github.com/Lightning-AI/pytorch-lightning/pull/20369))
- Allowed callbacks to be restored not just during training ([#20403](https://github.com/Lightning-AI/pytorch-lightning/pull/20403))
- Changed LightningCLI tests to account for future fix in jsonargparse ([#20372](https://github.com/Lightning-AI/pytorch-lightning/pull/20372))
- Bumped PyTorch to version `2.5` ([#20351](https://github.com/Lightning-AI/pytorch-lightning/pull/20351))
- Decoupled checkpoint artifact path from model artifact path ([#20325](https://github.com/Lightning-AI/pytorch-lightning/pull/20325))
- Updated BitsAndBytes version ([#20313](https://github.com/Lightning-AI/pytorch-lightning/pull/20313))
- Changed merging of hparams when logging to ignore parameter names that start with an underscore `_` ([#20221](https://github.com/Lightning-AI/pytorch-lightning/pull/20221))
- Re-enabled passing `BytesIO` as path in `.to_onnx()` ([#20172](https://github.com/Lightning-AI/pytorch-lightning/pull/20172))

### Removed

- Removed `List[int]` as input type for Trainer when `accelerator="cpu"` ([#20399](https://github.com/Lightning-AI/pytorch-lightning/pull/20399))

### Fixed

- Fixed UnboundLocalError when using the predict method with return_predictions=False. ([#20484](https://github.com/Lightning-AI/pytorch-lightning/pull/20484))
- Fixed use of `convert_module` in FSDP to avoid using more memory than necessary during initialization ([#20323](https://github.com/Lightning-AI/pytorch-lightning/pull/20323))
- Fixed TypeError in `configure_optimizers` when running with `ReduceLROnPlateau` ([#20471](https://github.com/Lightning-AI/pytorch-lightning/pull/20471))
- Fixed return type in `configure_optimizers` example ([#20420](https://github.com/Lightning-AI/pytorch-lightning/pull/20420))
- Fixed in ncorrect URI prefix stripping in MLFlowLogger ([#20365](https://github.com/Lightning-AI/pytorch-lightning/pull/20365))
- Fixed shuffling behavior when using a custom sampler in data module ([#20327](https://github.com/Lightning-AI/pytorch-lightning/pull/20327))
- Ensured restarting from checkpoints leads to consistent internal counters compared to uninterrupted training ([#20379](https://github.com/Lightnin…115548 tokens truncated…htning/pull/1349))
- Fixed checkpointing interval ([#1272](https://github.com/Lightning-AI/pytorch-lightning/pull/1272))
- Fixed validation and training loops run the partial dataset ([#1192](https://github.com/Lightning-AI/pytorch-lightning/pull/1192))
- Fixed running `on_validation_end` only on main process in DDP ([#1125](https://github.com/Lightning-AI/pytorch-lightning/pull/1125))
- Fixed `load_spawn_weights` only in proc rank 0 ([#1385](https://github.com/Lightning-AI/pytorch-lightning/pull/1385))
- Fixes using deprecated `use_amp` attribute ([#1145](https://github.com/Lightning-AI/pytorch-lightning/pull/1145))
- Fixed Tensorboard logger error: lightning_logs directory not exists in multi-node DDP on nodes with rank != 0 ([#1377](https://github.com/Lightning-AI/pytorch-lightning/pull/1377))
- Fixed `Unimplemented backend XLA` error on TPU ([#1387](https://github.com/Lightning-AI/pytorch-lightning/pull/1387))

## [0.7.1] - 2020-03-07

### Fixed

- Fixes `print` issues and `data_loader` ([#1080](https://github.com/Lightning-AI/pytorch-lightning/pull/1080))

## [0.7.0] - 2020-03-06

### Added

- Added automatic sampler setup. Depending on DDP or TPU, lightning configures the sampler correctly (user needs to do nothing) ([#926](https://github.com/Lightning-AI/pytorch-lightning/pull/926))
- Added `reload_dataloaders_every_epoch=False` flag for trainer. Some users require reloading data every epoch ([#926](https://github.com/Lightning-AI/pytorch-lightning/pull/926))
- Added `progress_bar_refresh_rate=50` flag for trainer. Throttle refresh rate on notebooks ([#926](https://github.com/Lightning-AI/pytorch-lightning/pull/926))
- Updated governance docs
- Added a check to ensure that the metric used for early stopping exists before training commences ([#542](https://github.com/Lightning-AI/pytorch-lightning/pull/542))
- Added `optimizer_idx` argument to `backward` hook ([#733](https://github.com/Lightning-AI/pytorch-lightning/pull/733))
- Added `entity` argument to `WandbLogger` to be passed to `wandb.init` ([#783](https://github.com/Lightning-AI/pytorch-lightning/pull/783))
- Added a tool for profiling training runs ([#782](https://github.com/Lightning-AI/pytorch-lightning/pull/782))
- Improved flexibility for naming of TensorBoard logs, can now set `version` to a `str` to just save to that directory, and use `name=''` to prevent experiment-name directory ([#804](https://github.com/Lightning-AI/pytorch-lightning/pull/804))
- Added option to specify `step` key when logging metrics ([#808](https://github.com/Lightning-AI/pytorch-lightning/pull/808))
- Added `train_dataloader`, `val_dataloader` and `test_dataloader` arguments to `Trainer.fit()`, for alternative data parsing ([#759](https://github.com/Lightning-AI/pytorch-lightning/pull/759))
- Added Tensor Processing Unit (TPU) support ([#868](https://github.com/Lightning-AI/pytorch-lightning/pull/868))
- Added semantic segmentation example ([#751](https://github.com/Lightning-AI/pytorch-lightning/pull/751),[#876](https://github.com/Lightning-AI/pytorch-lightning/pull/876),
     [#881](https://github.com/Lightning-AI/pytorch-lightning/pull/881))
- Split callbacks in multiple files ([#849](https://github.com/Lightning-AI/pytorch-lightning/pull/849))
- Support for user defined callbacks ([#889](https://github.com/Lightning-AI/pytorch-lightning/pull/889) and [#950](https://github.com/Lightning-AI/pytorch-lightning/pull/950))
- Added support for multiple loggers to be passed to `Trainer` as an iterable (e.g. list, tuple, etc.) ([#903](https://github.com/Lightning-AI/pytorch-lightning/pull/903))
- Added support for step-based learning rate scheduling ([#941](https://github.com/Lightning-AI/pytorch-lightning/pull/941))
- Added support for logging `hparams` as dict ([#1029](https://github.com/Lightning-AI/pytorch-lightning/pull/1029))
- Checkpoint and early stopping now work without val. step ([#1041](https://github.com/Lightning-AI/pytorch-lightning/pull/1041))
- Support graceful training cleanup after Keyboard Interrupt ([#856](https://github.com/Lightning-AI/pytorch-lightning/pull/856),
     [#1019](https://github.com/Lightning-AI/pytorch-lightning/pull/1019))
- Added type hints for function arguments ([#912](https://github.com/Lightning-AI/pytorch-lightning/pull/912), )
- Added default `argparser` for `Trainer` ([#952](https://github.com/Lightning-AI/pytorch-lightning/pull/1023),
     [#1023](https://github.com/Lightning-AI/pytorch-lightning/pull/1023))
- Added TPU gradient clipping ([#963](https://github.com/Lightning-AI/pytorch-lightning/pull/963))
- Added max/min number of steps in `Trainer` ([#728](https://github.com/Lightning-AI/pytorch-lightning/pull/728))

### Changed

- Improved `NeptuneLogger` by adding `close_after_fit` argument to allow logging after training([#908](https://github.com/Lightning-AI/pytorch-lightning/pull/1084))
- Changed default TQDM to use `tqdm.auto` for prettier outputs in IPython notebooks ([#752](https://github.com/Lightning-AI/pytorch-lightning/pull/752))
- Changed `pl.logging` to `pl.loggers` ([#767](https://github.com/Lightning-AI/pytorch-lightning/pull/767))
- Moved the default `tqdm_dict` definition from Trainer to `LightningModule`, so it can be overridden by the user ([#749](https://github.com/Lightning-AI/pytorch-lightning/pull/749))
- Moved functionality of `LightningModule.load_from_metrics` into `LightningModule.load_from_checkpoint` ([#995](https://github.com/Lightning-AI/pytorch-lightning/pull/995))
- Changed Checkpoint path parameter from `filepath` to `dirpath` ([#1016](https://github.com/Lightning-AI/pytorch-lightning/pull/1016))
- Freezed models `hparams` as `Namespace` property ([#1029](https://github.com/Lightning-AI/pytorch-lightning/pull/1029))
- Dropped `logging` config in package init ([#1015](https://github.com/Lightning-AI/pytorch-lightning/pull/1015))
- Renames model steps ([#1051](https://github.com/Lightning-AI/pytorch-lightning/pull/1051))
  - `training_end` >> `training_epoch_end`
  - `validation_end` >> `validation_epoch_end`
  - `test_end` >> `test_epoch_end`
- Refactor dataloading, supports infinite dataloader ([#955](https://github.com/Lightning-AI/pytorch-lightning/pull/955))
- Create single file in `TensorBoardLogger` ([#777](https://github.com/Lightning-AI/pytorch-lightning/pull/777))

### Deprecated

- Deprecated `pl.logging` ([#767](https://github.com/Lightning-AI/pytorch-lightning/pull/767))
- Deprecated `LightningModule.load_from_metrics` in favour of `LightningModule.load_from_checkpoint` ([#995](https://github.com/Lightning-AI/pytorch-lightning/pull/995),
     [#1079](https://github.com/Lightning-AI/pytorch-lightning/pull/1079))
- Deprecated `@data_loader` decorator ([#926](https://github.com/Lightning-AI/pytorch-lightning/pull/926))
- Deprecated model steps `training_end`, `validation_end` and `test_end` ([#1051](https://github.com/Lightning-AI/pytorch-lightning/pull/1051),
     [#1056](https://github.com/Lightning-AI/pytorch-lightning/pull/1056))

### Removed

- Removed dependency on `pandas` ([#736](https://github.com/Lightning-AI/pytorch-lightning/pull/736))
- Removed dependency on `torchvision` ([#797](https://github.com/Lightning-AI/pytorch-lightning/pull/797))
- Removed dependency on `scikit-learn` ([#801](https://github.com/Lightning-AI/pytorch-lightning/pull/801))

### Fixed

- Fixed a bug where early stopping `on_end_epoch` would be called inconsistently when `check_val_every_n_epoch == 0` ([#743](https://github.com/Lightning-AI/pytorch-lightning/pull/743))
- Fixed a bug where the model checkpointer didn't write to the same directory as the logger ([#771](https://github.com/Lightning-AI/pytorch-lightning/pull/771))
- Fixed a bug where the `TensorBoardLogger` class would create an additional empty log file during fitting ([#777](https://github.com/Lightning-AI/pytorch-lightning/pull/777))
- Fixed a bug where `global_step` was advanced incorrectly when using `accumulate_grad_batches > 1` ([#832](https://github.com/Lightning-AI/pytorch-lightning/pull/832))
- Fixed a bug when calling `self.logger.experiment` with multiple loggers ([#1009](https://github.com/Lightning-AI/pytorch-lightning/pull/1009))
- Fixed a bug when calling `logger.append_tags` on a `NeptuneLogger` with a single tag ([#1009](https://github.com/Lightning-AI/pytorch-lightning/pull/1009))
- Fixed sending back data from `.spawn` by saving and loading the trained model in/out of the process ([#1017](https://github.com/Lightning-AI/pytorch-lightning/pull/1017)
- Fixed port collision on DDP ([#1010](https://github.com/Lightning-AI/pytorch-lightning/pull/1010))
- Fixed/tested pass overrides ([#918](https://github.com/Lightning-AI/pytorch-lightning/pull/918))
- Fixed comet logger to log after train ([#892](https://github.com/Lightning-AI/pytorch-lightning/pull/892))
- Remove deprecated args to learning rate step function ([#890](https://github.com/Lightning-AI/pytorch-lightning/pull/890))

## [0.6.0] - 2020-01-21

### Added

- Added support for resuming from a specific checkpoint via `resume_from_checkpoint` argument ([#516](https://github.com/Lightning-AI/pytorch-lightning/pull/516))
- Added support for `ReduceLROnPlateau` scheduler ([#320](https://github.com/Lightning-AI/pytorch-lightning/pull/320))
- Added support for Apex mode `O2` in conjunction with Data Parallel ([#493](https://github.com/Lightning-AI/pytorch-lightning/pull/493))
- Added option (`save_top_k`) to save the top k models in the `ModelCheckpoint` class ([#128](https://github.com/Lightning-AI/pytorch-lightning/pull/128))
- Added `on_train_start` and `on_train_end` hooks to `ModelHooks` ([#598](https://github.com/Lightning-AI/pytorch-lightning/pull/598))
- Added `TensorBoardLogger` ([#607](https://github.com/Lightning-AI/pytorch-lightning/pull/607))
- Added support for weight summary of model with multiple inputs ([#543](https://github.com/Lightning-AI/pytorch-lightning/pull/543))
- Added `map_location` argument to `load_from_metrics` and `load_from_checkpoint` ([#625](https://github.com/Lightning-AI/pytorch-lightning/pull/625))
- Added option to disable validation by setting `val_percent_check=0` ([#649](https://github.com/Lightning-AI/pytorch-lightning/pull/649))
- Added `NeptuneLogger` class ([#648](https://github.com/Lightning-AI/pytorch-lightning/pull/648))
- Added `WandbLogger` class ([#627](https://github.com/Lightning-AI/pytorch-lightning/pull/627))

### Changed

- Changed the default progress bar to print to stdout instead of stderr ([#531](https://github.com/Lightning-AI/pytorch-lightning/pull/531))
- Renamed `step_idx` to `step`, `epoch_idx` to `epoch`, `max_num_epochs` to `max_epochs` and `min_num_epochs` to `min_epochs` ([#589](https://github.com/Lightning-AI/pytorch-lightning/pull/589))
- Renamed `total_batch_nb` to `total_batches`, `nb_val_batches` to `num_val_batches`, `nb_training_batches` to `num_training_batches`, `max_nb_epochs` to `max_epochs`, `min_nb_epochs` to `min_epochs`, `nb_test_batches` to `num_test_batches`, and `nb_val_batches` to `num_val_batches` ([#567](https://github.com/Lightning-AI/pytorch-lightning/pull/567))
- Changed gradient logging to use parameter names instead of indexes ([#660](https://github.com/Lightning-AI/pytorch-lightning/pull/660))
- Changed the default logger to `TensorBoardLogger` ([#609](https://github.com/Lightning-AI/pytorch-lightning/pull/609))
- Changed the directory for tensorboard logging to be the same as model checkpointing ([#706](https://github.com/Lightning-AI/pytorch-lightning/pull/706))

### Deprecated

- Deprecated `max_nb_epochs` and `min_nb_epochs` ([#567](https://github.com/Lightning-AI/pytorch-lightning/pull/567))
- Deprecated the `on_sanity_check_start` hook in `ModelHooks` ([#598](https://github.com/Lightning-AI/pytorch-lightning/pull/598))

### Removed

- Removed the `save_best_only` argument from `ModelCheckpoint`, use `save_top_k=1` instead ([#128](https://github.com/Lightning-AI/pytorch-lightning/pull/128))

### Fixed

- Fixed a bug which occurred when using Adagrad with cuda ([#554](https://github.com/Lightning-AI/pytorch-lightning/pull/554))
- Fixed a bug where training would be on the GPU despite setting `gpus=0` or `gpus=[]` ([#561](https://github.com/Lightning-AI/pytorch-lightning/pull/561))
- Fixed an error with `print_nan_gradients` when some parameters do not require gradient ([#579](https://github.com/Lightning-AI/pytorch-lightning/pull/579))
- Fixed a bug where the progress bar would show an incorrect number of total steps during the validation sanity check when using multiple validation data loaders ([#597](https://github.com/Lightning-AI/pytorch-lightning/pull/597))
- Fixed support for PyTorch 1.1.0 ([#552](https://github.com/Lightning-AI/pytorch-lightning/pull/552))
- Fixed an issue with early stopping when using a `val_check_interval < 1.0` in `Trainer` ([#492](https://github.com/Lightning-AI/pytorch-lightning/pull/492))
- Fixed bugs relating to the `CometLogger` object that would cause it to not work properly ([#481](https://github.com/Lightning-AI/pytorch-lightning/pull/481))
- Fixed a bug that would occur when returning `-1` from `on_batch_start` following an early exit or when the batch was `None` ([#509](https://github.com/Lightning-AI/pytorch-lightning/pull/509))
- Fixed a potential race condition with several processes trying to create checkpoint directories ([#530](https://github.com/Lightning-AI/pytorch-lightning/pull/530))
- Fixed a bug where batch 'segments' would remain on the GPU when using `truncated_bptt > 1` ([#532](https://github.com/Lightning-AI/pytorch-lightning/pull/532))
- Fixed a bug when using `IterableDataset` ([#547](https://github.com/Lightning-AI/pytorch-lightning/pull/547))
- Fixed a bug where `.item` was called on non-tensor objects ([#602](https://github.com/Lightning-AI/pytorch-lightning/pull/602))
- Fixed a bug where `Trainer.train` would crash on an uninitialized variable if the trainer was run after resuming from a checkpoint that was already at `max_epochs` ([#608](https://github.com/Lightning-AI/pytorch-lightning/pull/608))
- Fixed a bug where early stopping would begin two epochs early ([#617](https://github.com/Lightning-AI/pytorch-lightning/pull/617))
- Fixed a bug where `num_training_batches` and `num_test_batches` would sometimes be rounded down to zero ([#649](https://github.com/Lightning-AI/pytorch-lightning/pull/649))
- Fixed a bug where an additional batch would be processed when manually setting `num_training_batches` ([#653](https://github.com/Lightning-AI/pytorch-lightning/pull/653))
- Fixed a bug when batches did not have a `.copy` method ([#701](https://github.com/Lightning-AI/pytorch-lightning/pull/701))
- Fixed a bug when using `log_gpu_memory=True` in Python 3.6 ([#715](https://github.com/Lightning-AI/pytorch-lightning/pull/715))
- Fixed a bug where checkpoint writing could exit before completion, giving incomplete checkpoints ([#689](https://github.com/Lightning-AI/pytorch-lightning/pull/689))
- Fixed a bug where `on_train_end` was not called when ealy stopping ([#723](https://github.com/Lightning-AI/pytorch-lightning/pull/723))

## [0.5.3] - 2019-11-06

### Added

- Added option to disable default logger, checkpointer, and early stopping by passing `logger=False`, `checkpoint_callback=False` and `early_stop_callback=False` respectively
- Added `CometLogger` for use with Comet.ml
- Added `val_check_interval` argument to `Trainer` allowing validition to be performed at every given number of batches
- Added functionality to save and load hyperparameters using the standard checkpoint mechanism
- Added call to `torch.cuda.empty_cache` before training starts
- Added option for user to override the call t `backward`
- Added support for truncated backprop through time via the `truncated_bptt_steps` argument in `Trainer`
- Added option to operate on all outputs from `training_step` in DDP2
- Added a hook for modifying DDP init
- Added a hook for modifying Apex

### Changed

- Changed experiment version to be padded with zeros (e.g. `/dir/version_9` becomes `/dir/version_0009`)
- Changed callback metrics to include any metrics given in logs or progress bar
- Changed the default for `save_best_only` in `ModelCheckpoint` to `True`
- Added `tng_data_loader` for backwards compatibility
- Renamed `MLFlowLogger.client` to `MLFlowLogger.experiment` for consistency
- Moved `global_step` increment to happen after the batch has been processed
- Changed weights restore to first attempt HPC weights before restoring normally, preventing both weights being restored and running out of memory
- Changed progress bar functionality to add multiple progress bars for train/val/test
- Changed calls to `print` to use `logging` instead

### Deprecated

- Deprecated `tng_dataloader`

### Fixed

- Fixed an issue where the number of batches was off by one during training
- Fixed a bug that occurred when setting a checkpoint callback and `early_stop_callback=False`
- Fixed an error when importing CometLogger
- Fixed a bug where the `gpus` argument had some unexpected behaviour
- Fixed a bug where the computed total number of batches was sometimes incorrect
- Fixed a bug where the progress bar would sometimes not show the total number of batches in test mode
- Fixed a bug when using the `log_gpu_memory='min_max'` option in `Trainer`
- Fixed a bug where checkpointing would sometimes erase the current directory

## [0.5.2] - 2019-10-10

### Added

- Added `weights_summary` argument to `Trainer` to be set to `full` (full summary), `top` (just top level modules) or other
- Added `tags` argument to `MLFlowLogger`

### Changed

- Changed default for `amp_level` to `O1`

### Removed

- Removed the `print_weights_summary` argument from `Trainer`

### Fixed

- Fixed a bug where logs were not written properly
- Fixed a bug where `logger.finalize` wasn't called after training is complete
- Fixed callback metric errors in DDP
- Fixed a bug where `TestTubeLogger` didn't log to the correct directory

## [0.5.1] - 2019-10-05

### Added

- Added the `LightningLoggerBase` class for experiment loggers
- Added `MLFlowLogger` for logging with `mlflow`
- Added `TestTubeLogger` for logging with `test_tube`
- Added a different implementation of DDP (`distributed_backed='ddp2'`) where every node has one model using all GPUs
- Added support for optimisers which require a closure (e.g. LBFGS)
- Added automatic `MASTER_PORT` default for DDP when not set manually
- Added new GPU memory logging options `'min_max'` (log only the min/max utilization) and `'all'` (log all the GPU memory)

### Changed

- Changed schedulers to always be called with the current epoch
- Changed `test_tube` to an optional dependency
- Changed data loaders to internally use a getter instead of a python property
- Disabled auto GPU loading when restoring weights to prevent out of memory errors
- Changed logging, early stopping and checkpointing to occur by default

### Fixed

- Fixed a bug with samplers that do not specify `set_epoch`
- Fixed a bug when using the `MLFlowLogger` with unsupported data types, this will now raise a warning
- Fixed a bug where gradient norms were always zero using `track_grad_norm`
- Fixed a bug which causes a crash when logging memory

## [0.5.0] - 2019-09-26

### Changed

- Changed `data_batch` argument to `batch` throughout
- Changed `batch_i` argument to `batch_idx` throughout
- Changed `tng_dataloader` method to `train_dataloader`
- Changed `on_tng_metrics` method to `on_training_metrics`
- Changed `gradient_clip` argument to `gradient_clip_val`
- Changed `add_log_row_interval` to `row_log_interval`

### Fixed

- Fixed a bug with tensorboard logging in multi-gpu setup

## [0.4.9] - 2019-09-16

### Added

- Added the flag `log_gpu_memory` to `Trainer` to deactivate logging of GPU memory utilization
- Added SLURM resubmit functionality (port from test-tube)
- Added optional weight_save_path to trainer to remove the need for a checkpoint_callback when using cluster training
- Added option to use single gpu per node with `DistributedDataParallel`

### Changed

- Changed functionality of `validation_end` and `test_end` with multiple dataloaders to be given all of the dataloaders at once rather than in separate calls
- Changed print_nan_grads to only print the parameter value and gradients when they contain NaN
- Changed gpu API to take integers as well (e.g. `gpus=2` instead of `gpus=[0, 1]`)
- All models now loaded on to CPU to avoid device and out of memory issues in PyTorch

### Fixed

- Fixed a bug where data types that implement `.to` but not `.cuda` would not be properly moved onto the GPU
- Fixed a bug where data would not be re-shuffled every epoch when using a `DistributedSampler`

## [0.4.8] - 2019-08-31

### Added

- Added `test_step` and `test_end` methods, used when `Trainer.test` is called
- Added `GradientAccumulationScheduler` callback which can be used to schedule changes to the number of accumulation batches
- Added option to skip the validation sanity check by setting `nb_sanity_val_steps = 0`

### Fixed

- Fixed a bug when setting `nb_sanity_val_steps = 0`

## [0.4.7] - 2019-08-24

### Changed

- Changed the default `val_check_interval` to `1.0`
- Changed defaults for `nb_val_batches`, `nb_tng_batches` and `nb_test_batches` to 0

### Fixed

- Fixed a bug where the full validation set as used despite setting `val_percent_check`
- Fixed a bug where an `Exception` was thrown when using a data set containing a single batch
- Fixed a bug where an `Exception` was thrown if no `val_dataloader` was given
- Fixed a bug where tuples were not properly transferred to the GPU
- Fixed a bug where data of a non standard type was not properly handled by the trainer
- Fixed a bug when loading data as a tuple
- Fixed a bug where `AttributeError` could be suppressed by the `Trainer`

## [0.4.6] - 2019-08-15

### Added

- Added support for data to be given as a `dict` or `list` with a single gpu
- Added support for `configure_optimizers` to return a single optimizer, two list (optimizers and schedulers), or a single list

### Fixed

- Fixed a bug where returning just an optimizer list (i.e. without schedulers) from `configure_optimizers` would throw an `Exception`

## [0.4.5] - 2019-08-13

### Added

- Added `optimizer_step` method that can be overridden to change the standard optimizer behaviour

## [0.4.4] - 2019-08-12

### Added

- Added support for multiple validation dataloaders
- Added support for latest test-tube logger (optimised for `torch==1.2.0`)

### Changed

- `validation_step` and `val_dataloader` are now optional
- `lr_scheduler` is now activated after epoch

### Fixed

- Fixed a bug where a warning would show when using `lr_scheduler` in `torch>1.1.0`
- Fixed a bug where an `Exception` would be thrown if using `torch.DistributedDataParallel` without using a `DistributedSampler`, this now throws a `Warning` instead

## [0.4.3] - 2019-08-10

### Fixed

- Fixed a bug where accumulate gradients would scale the loss incorrectly

## [0.4.2] - 2019-08-08

### Changed

- Changed install requirement to `torch==1.2.0`

## [0.4.1] - 2019-08-08

### Changed

- Changed install requirement to `torch==1.1.0`

## [0.4.0] - 2019-08-08

### Added

- Added 16-bit support for a single GPU
- Added support for training continuation (preserves epoch, global step etc.)

### Changed

- Changed `training_step` and `validation_step`, outputs will no longer be automatically reduced

### Removed

- Removed need for `Experiment` object in `Trainer`

### Fixed

- Fixed issues with reducing outputs from generative models (such as images and text)

## [0.3.6] - 2019-07-25

### Added

- Added a decorator to do lazy data loading internally

### Fixed

- Fixed a bug where `Experiment` object was not process safe, potentially causing logs to be overwritten

## [0.3.5] - 2019-07-25

## [0.3.4] - 2019-07-22

## [0.3.3] - 2019-07-22

## [0.3.2] - 2019-07-21

## [0.3.1] - 2019-07-21

## [0.2.x] - 2019-07-09

## [0.1.x] - 2019-06-DD
